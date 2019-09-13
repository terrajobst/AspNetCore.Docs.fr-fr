---
title: Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core
author: guardrex
description: Obtenez des conseils de résolution de problèmes pour les erreurs courantes liées à l’hébergement d’applications ASP.NET Core sur Azure Apps Service et IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: f6afd6491181830f4d79486fa26a64423cd4a0ac
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963672"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="19f72-103">Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19f72-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="19f72-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="19f72-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="19f72-105">Cette rubrique décrit les erreurs courantes et fournit des conseils de dépannage pour les erreurs spécifiques lors de l’hébergement d’applications ASP.NET Core sur Azure Apps service et IIS.</span><span class="sxs-lookup"><span data-stu-id="19f72-105">This topic describes common errors and provides troubleshooting advice for specific errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="19f72-106">Pour obtenir des instructions générales sur <xref:test/troubleshoot-azure-iis>la résolution des problèmes, consultez.</span><span class="sxs-lookup"><span data-stu-id="19f72-106">For general troubleshooting guidance, see <xref:test/troubleshoot-azure-iis>.</span></span>

<span data-ttu-id="19f72-107">Collectez les informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="19f72-107">Collect the following information:</span></span>

* <span data-ttu-id="19f72-108">Comportement du navigateur (code d’état et message d’erreur)</span><span class="sxs-lookup"><span data-stu-id="19f72-108">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="19f72-109">Entrées du journal des événements de l’application</span><span class="sxs-lookup"><span data-stu-id="19f72-109">Application Event Log entries</span></span>
  * <span data-ttu-id="19f72-110">Azure App Service &ndash; Consultez <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="19f72-110">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="19f72-111">IIS</span><span class="sxs-lookup"><span data-stu-id="19f72-111">IIS</span></span>
    1. <span data-ttu-id="19f72-112">Sélectionnez **Démarrer** dans le menu **Windows**, tapez *Observateur d’événements*, puis appuyez sur **Entrée**.</span><span class="sxs-lookup"><span data-stu-id="19f72-112">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="19f72-113">Une fois l’**Observateur d’événements** ouvert, développez **Journaux Windows** > **Application** dans la barre latérale.</span><span class="sxs-lookup"><span data-stu-id="19f72-113">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="19f72-114">Entrées de journal stdout et de débogage du module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19f72-114">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="19f72-115">Azure App Service &ndash; Consultez <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="19f72-115">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="19f72-116">IIS &ndash; Suivez les instructions données dans les sections [Création et redirection de journal](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) et [Journaux de diagnostic améliorés](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) de la rubrique Module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="19f72-116">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="19f72-117">Comparez les informations d’erreur aux erreurs courantes suivantes.</span><span class="sxs-lookup"><span data-stu-id="19f72-117">Compare error information to the following common errors.</span></span> <span data-ttu-id="19f72-118">Si vous trouvez une correspondance, suivez les conseils de dépannage.</span><span class="sxs-lookup"><span data-stu-id="19f72-118">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="19f72-119">La liste d’erreurs de cette rubrique n’est pas exhaustive.</span><span class="sxs-lookup"><span data-stu-id="19f72-119">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="19f72-120">Si vous rencontrez une erreur non listée ici, ouvrez un nouveau problème à l’aide du bouton de **Commentaires sur le contenu** situé au bas de cette rubrique, puis fournissez des instructions détaillées sur la manière de reproduire l’erreur.</span><span class="sxs-lookup"><span data-stu-id="19f72-120">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="19f72-121">Le programme d’installation ne parvient pas à obtenir VC++ Redistributable</span><span class="sxs-lookup"><span data-stu-id="19f72-121">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="19f72-122">**Exception du programme d’installation :** 0x80072efd **--OU--** 0x80072f76 - Erreur non spécifiée</span><span class="sxs-lookup"><span data-stu-id="19f72-122">**Installer Exception:** 0x80072efd **--OR--** 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="19f72-123">**Exception du journal d’installation&#8224; :** Erreur 0x80072efd **--OU--** 0x80072f76 : Échec de l’exécution du package EXE</span><span class="sxs-lookup"><span data-stu-id="19f72-123">**Installer Log Exception&#8224;:** Error 0x80072efd **--OR--** 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="19f72-124">&#8224;Le journal se trouve sur *C:\Users\{UTILISATEUR}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{HORODATAGE}.log*.</span><span class="sxs-lookup"><span data-stu-id="19f72-124">&#8224;The log is located at *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span></span>

<span data-ttu-id="19f72-125">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="19f72-125">Troubleshooting:</span></span>

<span data-ttu-id="19f72-126">Si le système n’a pas accès à Internet au moment de l’[installation du bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), cette exception se produit quand le programme d’installation ne peut pas obtenir *Microsoft Visual C++ 2015 Redistributable*.</span><span class="sxs-lookup"><span data-stu-id="19f72-126">If the system doesn't have Internet access while [installing the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="19f72-127">Obtenez un programme d’installation à partir du [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="19f72-127">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="19f72-128">En cas d’échec du programme d’installation, le serveur risque de ne pas recevoir le runtime .NET Core nécessaire à l’hébergement d’un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="19f72-128">If the installer fails, the server may not receive the .NET Core runtime required to host a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span> <span data-ttu-id="19f72-129">En cas d’hébergement d’un déploiement dépendant du framework, vérifiez que le runtime est installé dans **Programmes et fonctionnalités** ou **Applications et fonctionnalités**.</span><span class="sxs-lookup"><span data-stu-id="19f72-129">If hosting an FDD, confirm that the runtime is installed in **Programs & Features** or **Apps & features**.</span></span> <span data-ttu-id="19f72-130">Si un runtime spécifique est nécessaire, téléchargez-le à partir des [archives de téléchargement .NET](https://dotnet.microsoft.com/download/archives), puis installez-le sur le système.</span><span class="sxs-lookup"><span data-stu-id="19f72-130">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="19f72-131">Après avoir installé le runtime, redémarrez le système ou IIS en exécutant **net stop was /y** suivi de **net start w3svc** à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="19f72-131">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="19f72-132">La mise à niveau du système d’exploitation a supprimé le Module ASP.NET Core 32 bits</span><span class="sxs-lookup"><span data-stu-id="19f72-132">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="19f72-133">**Journal des applications :** Le fichier DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** du module n’a pas pu se charger.</span><span class="sxs-lookup"><span data-stu-id="19f72-133">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="19f72-134">Les données sont erronées.</span><span class="sxs-lookup"><span data-stu-id="19f72-134">The data is the error.</span></span>

<span data-ttu-id="19f72-135">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="19f72-135">Troubleshooting:</span></span>

<span data-ttu-id="19f72-136">Les fichiers autres que les fichiers de système d’exploitation dans le répertoire **C:\Windows\SysWOW64\inetsrv** ne sont pas conservés pendant la mise à niveau du système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="19f72-136">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="19f72-137">Si le module ASP.NET Core est installé avant la mise à niveau d’un système d’exploitation et si un pool d’applications est exécuté en mode 32 bits après la mise à niveau du système d’exploitation, ce problème se produit.</span><span class="sxs-lookup"><span data-stu-id="19f72-137">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="19f72-138">Après une mise à niveau du système d’exploitation, réparez le Module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="19f72-138">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="19f72-139">Consultez [Installer le bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="19f72-139">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="19f72-140">Sélectionnez **Réparer** quand le programme d’installation est exécuté.</span><span class="sxs-lookup"><span data-stu-id="19f72-140">Select **Repair** when the installer is run.</span></span>

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a><span data-ttu-id="19f72-141">Extension de site manquante, extensions de site 32 bits (x86) et 64 bits (x64) installées ou nombre de bits de processus incorrect défini</span><span class="sxs-lookup"><span data-stu-id="19f72-141">Missing site extension, 32-bit (x86) and 64-bit (x64) site extensions installed, or wrong process bitness set</span></span>

<span data-ttu-id="19f72-142">*S’applique aux applications hébergées par Azure App Services.*</span><span class="sxs-lookup"><span data-stu-id="19f72-142">*Applies to apps hosted by Azure App Services.*</span></span>

* <span data-ttu-id="19f72-143">**Navigateur :** Erreur HTTP 500.0 - Échec du chargement du gestionnaire in-process d’ANCM</span><span class="sxs-lookup"><span data-stu-id="19f72-143">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="19f72-144">**Journal des applications :** Échec de l’appel de hostfxr pour la recherche du gestionnaire de requêtes in-process. Dépendances natives introuvables.</span><span class="sxs-lookup"><span data-stu-id="19f72-144">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="19f72-145">Le gestionnaire de requêtes in-process est introuvable.</span><span class="sxs-lookup"><span data-stu-id="19f72-145">Could not find inprocess request handler.</span></span> <span data-ttu-id="19f72-146">Sortie capturée à partir de l’appel de hostfxr : Il n’a pas été possible de localiser une version compatible du framework.</span><span class="sxs-lookup"><span data-stu-id="19f72-146">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="19f72-147">Le framework spécifié « Microsoft.AspNetCore.App », version « {VERSION}-preview-\* » est introuvable.</span><span class="sxs-lookup"><span data-stu-id="19f72-147">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span> <span data-ttu-id="19f72-148">Échec du démarrage de l’application « /LM/W3SVC/1416782824/ROOT ». Code d’erreur : 0x8000ffff.</span><span class="sxs-lookup"><span data-stu-id="19f72-148">Failed to start application '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="19f72-149">**Journal stdout du module ASP.NET Core :** Il n’a pas été possible de localiser une version compatible du framework.</span><span class="sxs-lookup"><span data-stu-id="19f72-149">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="19f72-150">Le framework spécifié « Microsoft.AspNetCore.App », version « {VERSION}-preview-\* » est introuvable.</span><span class="sxs-lookup"><span data-stu-id="19f72-150">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="19f72-151">**Journal de débogage du module ASP.NET Core :** Échec de l’appel de hostfxr pour la recherche du gestionnaire de requêtes in-process. Dépendances natives introuvables.</span><span class="sxs-lookup"><span data-stu-id="19f72-151">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="19f72-152">Cela signifie très probablement que l’application est mal configurée. Vérifiez les versions de Microsoft.NetCore.App et Microsoft.AspNetCore.App ciblées par l’application et installées sur la machine.</span><span class="sxs-lookup"><span data-stu-id="19f72-152">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="19f72-153">HRESULT incorrect retourné : 0x8000ffff.</span><span class="sxs-lookup"><span data-stu-id="19f72-153">Failed HRESULT returned: 0x8000ffff.</span></span> <span data-ttu-id="19f72-154">Le gestionnaire de requêtes in-process est introuvable.</span><span class="sxs-lookup"><span data-stu-id="19f72-154">Could not find inprocess request handler.</span></span> <span data-ttu-id="19f72-155">Il n’a pas été possible de localiser une version compatible du framework.</span><span class="sxs-lookup"><span data-stu-id="19f72-155">It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="19f72-156">Le framework spécifié « Microsoft.AspNetCore.App », version « {VERSION}-preview-\* » est introuvable.</span><span class="sxs-lookup"><span data-stu-id="19f72-156">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker-end

<span data-ttu-id="19f72-157">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="19f72-157">Troubleshooting:</span></span>

* <span data-ttu-id="19f72-158">Si vous exécutez l’application sur un runtime en préversion, installez l’extension de site 32 bits (x86) **ou** 64 bits (x64) qui correspond au nombre de bits de l’application et à la version du runtime de l’application.</span><span class="sxs-lookup"><span data-stu-id="19f72-158">If running the app on a preview runtime, install either the 32-bit (x86) **or** 64-bit (x64) site extension that matches the bitness of the app and the app's runtime version.</span></span> <span data-ttu-id="19f72-159">**N’installez pas les deux extensions ou plusieurs versions du runtime de l’extension.**</span><span class="sxs-lookup"><span data-stu-id="19f72-159">**Don't install both extensions or multiple runtime versions of the extension.**</span></span>

  * <span data-ttu-id="19f72-160">Runtime ASP.NET Core {RUNTIME VERSION} (x86)</span><span class="sxs-lookup"><span data-stu-id="19f72-160">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span></span>
  * <span data-ttu-id="19f72-161">Runtime ASP.NET Core {RUNTIME VERSION} (x64)</span><span class="sxs-lookup"><span data-stu-id="19f72-161">ASP.NET Core {RUNTIME VERSION} (x64) Runtime</span></span>

  <span data-ttu-id="19f72-162">Redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="19f72-162">Restart the app.</span></span> <span data-ttu-id="19f72-163">Patientez quelques secondes pour que l’application redémarre.</span><span class="sxs-lookup"><span data-stu-id="19f72-163">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="19f72-164">Si vous exécutez l’application sur un runtime en préversion et que les deux [extensions de site](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) 32 bits (x86) et 64 bits (x64) sont installées, désinstallez l’extension de site qui ne correspond pas au nombre de bits de l’application.</span><span class="sxs-lookup"><span data-stu-id="19f72-164">If running the app on a preview runtime and both the 32-bit (x86) and 64-bit (x64) [site extensions](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) are installed, uninstall the site extension that doesn't match the bitness of the app.</span></span> <span data-ttu-id="19f72-165">Après avoir supprimé l’extension de site, redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="19f72-165">After removing the site extension, restart the app.</span></span> <span data-ttu-id="19f72-166">Patientez quelques secondes pour que l’application redémarre.</span><span class="sxs-lookup"><span data-stu-id="19f72-166">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="19f72-167">Si vous exécutez l’application sur un runtime en préversion et que le nombre de bits de l’extension de site correspond à celui de l’application, vérifiez que la *version du runtime* de l’extension de site en préversion correspond à la version du runtime de l’application.</span><span class="sxs-lookup"><span data-stu-id="19f72-167">If running the app on a preview runtime and the site extension's bitness matches that of the app, confirm that the preview site extension's *runtime version* matches the app's runtime version.</span></span>

* <span data-ttu-id="19f72-168">Vérifiez que la **plateforme** de l’application dans **Paramètres de l’application** correspond au nombre de bits de l’application.</span><span class="sxs-lookup"><span data-stu-id="19f72-168">Confirm that the app's **Platform** in **Application Settings** matches the bitness of the app.</span></span>

<span data-ttu-id="19f72-169">Pour plus d'informations, consultez <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span><span class="sxs-lookup"><span data-stu-id="19f72-169">For more information, see <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="19f72-170">Une application x86 est déployée mais le pool d’applications n’est pas activé pour les applications 32 bits</span><span class="sxs-lookup"><span data-stu-id="19f72-170">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="19f72-171">**Navigateur :** Erreur HTTP 500.30 - Échec du démarrage in-process d’ANCM</span><span class="sxs-lookup"><span data-stu-id="19f72-171">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="19f72-172">**Journal des applications :** L’application « /LM/W3SVC/5/ROOT » ayant pour racine physique « {PATH} » a rencontré une exception managée inattendue. Code d’exception = « 0xe0434352 ».</span><span class="sxs-lookup"><span data-stu-id="19f72-172">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="19f72-173">Pour plus d’informations, consultez les journaux stderr.</span><span class="sxs-lookup"><span data-stu-id="19f72-173">Please check the stderr logs for more information.</span></span> <span data-ttu-id="19f72-174">L’application « /LM/W3SVC/5/ROOT » ayant pour racine physique « {PATH} » n’a pas pu charger clr et l’application managée.</span><span class="sxs-lookup"><span data-stu-id="19f72-174">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="19f72-175">Sortie prématurée du thread de travail CLR</span><span class="sxs-lookup"><span data-stu-id="19f72-175">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="19f72-176">**Journal stdout du module ASP.NET Core :** Le fichier journal est créé mais il est vide.</span><span class="sxs-lookup"><span data-stu-id="19f72-176">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="19f72-177">**Journal de débogage du module ASP.NET Core :** HRESULT incorrect retourné : 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="19f72-177">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

<span data-ttu-id="19f72-178">Ce scénario est intercepté par le kit SDK au moment de la publication d’une application autonome.</span><span class="sxs-lookup"><span data-stu-id="19f72-178">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="19f72-179">Le kit SDK génère une erreur si le RID ne correspond pas à la cible de la plateforme (par exemple, un RID `win10-x64` avec `<PlatformTarget>x86</PlatformTarget>` dans le fichier projet).</span><span class="sxs-lookup"><span data-stu-id="19f72-179">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="19f72-180">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="19f72-180">Troubleshooting:</span></span>

<span data-ttu-id="19f72-181">Pour un déploiement dépendant du framework x86 (`<PlatformTarget>x86</PlatformTarget>`), activez le pool d’applications IIS pour les applications 32 bits.</span><span class="sxs-lookup"><span data-stu-id="19f72-181">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="19f72-182">Dans le Gestionnaire IIS, ouvrez les **Paramètres avancés** du pool d’applications, puis affectez à l’option **Activer les applications 32 bits** la valeur **Vrai**.</span><span class="sxs-lookup"><span data-stu-id="19f72-182">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="19f72-183">Conflits de plateforme avec RID</span><span class="sxs-lookup"><span data-stu-id="19f72-183">Platform conflicts with RID</span></span>

* <span data-ttu-id="19f72-184">**Navigateur :** Erreur HTTP 502.5 - Échec du processus</span><span class="sxs-lookup"><span data-stu-id="19f72-184">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="19f72-185">**Journal des applications :** L’application « MACHINE/WEBROOT/APPHOST/{ASSEMBLY} » ayant pour racine physique « C:\{CHEMIN}\' » n’a pas pu démarrer le processus à l’aide de la ligne de commande « C:\{CHEMIN}{ASSEMBLY}.{exe|dll} ». Code d’erreur = « 0x80004005 : ff ».</span><span class="sxs-lookup"><span data-stu-id="19f72-185">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="19f72-186">**Journal stdout du module ASP.NET Core :** Exception non prise en charge : System.BadImageFormatException : Impossible de charger le fichier ou l’assembly « {ASSEMBLY}.dll ».</span><span class="sxs-lookup"><span data-stu-id="19f72-186">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="19f72-187">Tentative de chargement d’un programme au format incorrect.</span><span class="sxs-lookup"><span data-stu-id="19f72-187">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="19f72-188">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="19f72-188">Troubleshooting:</span></span>

* <span data-ttu-id="19f72-189">Vérifiez que l’application s’exécute localement sur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="19f72-189">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="19f72-190">Un échec de processus peut être dû à un problème au niveau de l’application.</span><span class="sxs-lookup"><span data-stu-id="19f72-190">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="19f72-191">Pour plus d'informations, consultez <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="19f72-191">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="19f72-192">Si cette exception se produit pour un déploiement d’applications Azure pendant la mise à niveau d’une application et le déploiement de nouveaux assemblys, supprimez manuellement tous les fichiers du déploiement précédent.</span><span class="sxs-lookup"><span data-stu-id="19f72-192">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="19f72-193">Le fait de laisser des assemblys incompatibles peut provoquer une exception `System.BadImageFormatException` lors du déploiement d’une application mise à niveau.</span><span class="sxs-lookup"><span data-stu-id="19f72-193">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="19f72-194">Point de terminaison d’URI incorrect ou site web arrêté</span><span class="sxs-lookup"><span data-stu-id="19f72-194">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="19f72-195">**Navigateur :** ERR_CONNECTION_REFUSED **--OU--** Connexion impossible</span><span class="sxs-lookup"><span data-stu-id="19f72-195">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="19f72-196">**Journal des applications :** Entrée interdite</span><span class="sxs-lookup"><span data-stu-id="19f72-196">**Application Log:** No entry</span></span>

* <span data-ttu-id="19f72-197">**Journal stdout du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="19f72-197">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="19f72-198">**Journal de débogage du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="19f72-198">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="19f72-199">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="19f72-199">Troubleshooting:</span></span>

* <span data-ttu-id="19f72-200">Vérifiez que le point de terminaison d’URI approprié de l’application est en cours d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="19f72-200">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="19f72-201">Vérifiez les liaisons.</span><span class="sxs-lookup"><span data-stu-id="19f72-201">Check the bindings.</span></span>

* <span data-ttu-id="19f72-202">Vérifiez que le site web IIS n’est pas à l’état *Arrêté*.</span><span class="sxs-lookup"><span data-stu-id="19f72-202">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="19f72-203">Fonctionnalités de serveur W3SVC ou CoreWebEngine désactivées</span><span class="sxs-lookup"><span data-stu-id="19f72-203">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="19f72-204">**Exception de système d’exploitation :** Les fonctionnalités CoreWebEngine et W3SVC d’IIS 7.0 doivent être installées pour permettre l’utilisation du module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="19f72-204">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="19f72-205">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="19f72-205">Troubleshooting:</span></span>

<span data-ttu-id="19f72-206">Vérifiez que le rôle et les fonctionnalités appropriés sont activés.</span><span class="sxs-lookup"><span data-stu-id="19f72-206">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="19f72-207">Consultez [Configuration d’IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="19f72-207">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="19f72-208">Chemin physique de site web incorrect ou application manquante</span><span class="sxs-lookup"><span data-stu-id="19f72-208">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="19f72-209">**Navigateur :** 403 Interdit - Accès refusé **--OU--** 403.14 Interdit - Le serveur web est configuré pour ne pas afficher le contenu de ce répertoire.</span><span class="sxs-lookup"><span data-stu-id="19f72-209">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="19f72-210">**Journal des applications :** Entrée interdite</span><span class="sxs-lookup"><span data-stu-id="19f72-210">**Application Log:** No entry</span></span>

* <span data-ttu-id="19f72-211">**Journal stdout du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="19f72-211">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="19f72-212">**Journal de débogage du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="19f72-212">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="19f72-213">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="19f72-213">Troubleshooting:</span></span>

<span data-ttu-id="19f72-214">Consultez les **Paramètres de base** du site web IIS et le dossier d’application physique.</span><span class="sxs-lookup"><span data-stu-id="19f72-214">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="19f72-215">Vérifiez que l’application est dans le dossier sur le **chemin physique** du site web IIS.</span><span class="sxs-lookup"><span data-stu-id="19f72-215">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="19f72-216">Rôle incorrect, module ASP.NET Core non installé ou autorisations incorrectes</span><span class="sxs-lookup"><span data-stu-id="19f72-216">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="19f72-217">**Navigateur :** 500.19 Erreur de serveur interne - Impossible d’accéder à la page demandée, car les données de configuration associées à la page sont non valides.</span><span class="sxs-lookup"><span data-stu-id="19f72-217">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="19f72-218">**--OU--** Impossible d’afficher cette page</span><span class="sxs-lookup"><span data-stu-id="19f72-218">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="19f72-219">**Journal des applications :** Entrée interdite</span><span class="sxs-lookup"><span data-stu-id="19f72-219">**Application Log:** No entry</span></span>

* <span data-ttu-id="19f72-220">**Journal stdout du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="19f72-220">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="19f72-221">**Journal de débogage du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="19f72-221">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="19f72-222">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="19f72-222">Troubleshooting:</span></span>

* <span data-ttu-id="19f72-223">Vérifiez que le rôle approprié est activé.</span><span class="sxs-lookup"><span data-stu-id="19f72-223">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="19f72-224">Consultez [Configuration d’IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="19f72-224">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="19f72-225">Ouvrez **Programmes et fonctionnalités** ou **Applications et fonctionnalités**, puis vérifiez que **Windows Server Hosting** est installé.</span><span class="sxs-lookup"><span data-stu-id="19f72-225">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="19f72-226">Si **Windows Server Hosting** ne figure pas dans la liste des programmes installés, téléchargez et installez le bundle d’hébergement .NET Core.</span><span class="sxs-lookup"><span data-stu-id="19f72-226">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="19f72-227">Programme d’installation du bundle d’hébergement .NET Core actuel (téléchargement direct)</span><span class="sxs-lookup"><span data-stu-id="19f72-227">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="19f72-228">Pour plus d’informations, consultez [Installer le bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="19f72-228">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="19f72-229">Vérifiez que **Pool d’applications** > **Modèle de processus** > **Identité** a la valeur **ApplicationPoolIdentity** ou que l’identité personnalisée dispose des autorisations appropriées pour accéder au dossier de déploiement de l’application.</span><span class="sxs-lookup"><span data-stu-id="19f72-229">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="19f72-230">Si vous avez désinstallé le bundle d’hébergement ASP.NET Core et installé une version antérieure du bundle d’hébergement, le fichier *applicationHost.config* ne contient pas de section pour le module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="19f72-230">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="19f72-231">Ouvrez *applicationHost.config* sur *%windir%/System32/inetsrv/config* et recherchez le groupe de sections `<configuration><configSections><sectionGroup name="system.webServer">`.</span><span class="sxs-lookup"><span data-stu-id="19f72-231">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="19f72-232">Si la section pour le module ASP.NET Core ne se trouve pas dans le groupe de sections, ajoutez l’élément de section :</span><span class="sxs-lookup"><span data-stu-id="19f72-232">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  <span data-ttu-id="19f72-233">Sinon, installez la dernière version du bundle d’hébergement ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="19f72-233">Alternatively, install the latest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="19f72-234">La dernière version est rétrocompatible avec les applications ASP.NET Core prises en charge.</span><span class="sxs-lookup"><span data-stu-id="19f72-234">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="19f72-235">processPath incorrect, variable de chemin manquante, bundle d’hébergement non installé, système/IIS non redémarré, VC++ Redistributable non installé ou violation d’accès dotnet.exe</span><span class="sxs-lookup"><span data-stu-id="19f72-235">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="19f72-236">**Navigateur :** Erreur HTTP 500.0 - Échec du chargement du gestionnaire in-process d’ANCM</span><span class="sxs-lookup"><span data-stu-id="19f72-236">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="19f72-237">**Journal des applications :** L’application « MACHINE/WEBROOT/APPHOST/{ASSEMBLY} » ayant pour racine physique « C:\{CHEMIN}\' » n’a pas pu démarrer le processus à l’aide de la ligne de commande « {...} »</span><span class="sxs-lookup"><span data-stu-id="19f72-237">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="19f72-238">Code d’erreur = 0x80070002 : 0.</span><span class="sxs-lookup"><span data-stu-id="19f72-238">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="19f72-239">L’application « {PATH} » n’a pas pu démarrer.</span><span class="sxs-lookup"><span data-stu-id="19f72-239">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="19f72-240">L’exécutable est introuvable sur « {PATH} ».</span><span class="sxs-lookup"><span data-stu-id="19f72-240">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="19f72-241">Échec du démarrage de l’application « /LM/W3SVC/2/ROOT ». Code d’erreur : 0x8007023e.</span><span class="sxs-lookup"><span data-stu-id="19f72-241">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="19f72-242">**Journal stdout du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="19f72-242">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="19f72-243">**Journal de débogage du module ASP.NET Core :** Journal des événements : L’application « {PATH} » n’a pas pu démarrer.</span><span class="sxs-lookup"><span data-stu-id="19f72-243">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="19f72-244">L’exécutable est introuvable sur « {PATH} ».</span><span class="sxs-lookup"><span data-stu-id="19f72-244">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="19f72-245">HRESULT incorrect retourné : 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="19f72-245">Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="19f72-246">**Navigateur :** Erreur HTTP 502.5 - Échec du processus</span><span class="sxs-lookup"><span data-stu-id="19f72-246">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="19f72-247">**Journal des applications :** L’application « MACHINE/WEBROOT/APPHOST/{ASSEMBLY} » ayant pour racine physique « C:\{CHEMIN}\' » n’a pas pu démarrer le processus à l’aide de la ligne de commande « {...} »</span><span class="sxs-lookup"><span data-stu-id="19f72-247">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="19f72-248">Code d’erreur = 0x80070002 : 0.</span><span class="sxs-lookup"><span data-stu-id="19f72-248">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="19f72-249">**Journal stdout du module ASP.NET Core :** Le fichier journal est créé mais il est vide.</span><span class="sxs-lookup"><span data-stu-id="19f72-249">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="19f72-250">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="19f72-250">Troubleshooting:</span></span>

* <span data-ttu-id="19f72-251">Vérifiez que l’application s’exécute localement sur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="19f72-251">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="19f72-252">Un échec de processus peut être dû à un problème au niveau de l’application.</span><span class="sxs-lookup"><span data-stu-id="19f72-252">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="19f72-253">Pour plus d'informations, consultez <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="19f72-253">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="19f72-254">Examinez l’attribut *processPath* de l’élément `<aspNetCore>` dans *web.config* afin de vérifier qu’il s’agit de `dotnet` pour un déploiement dépendant du framework ou de `.\{ASSEMBLY}.exe` pour un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="19f72-254">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="19f72-255">Pour un déploiement dépendant du framework, *dotnet.exe* peut ne pas être accessible via les paramètres PATH.</span><span class="sxs-lookup"><span data-stu-id="19f72-255">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="19f72-256">Vérifiez que *C:\Program Files\dotnet\\* existe dans les paramètres PATH du système.</span><span class="sxs-lookup"><span data-stu-id="19f72-256">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="19f72-257">Dans le cas d’un déploiement dépendant du framework, *dotnet.exe* risque de ne pas être accessible pour l’identité de l’utilisateur du pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="19f72-257">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="19f72-258">Vérifiez que l’identité de l’utilisateur du pool d’applications a accès au répertoire *C:\Program Files\dotnet*.</span><span class="sxs-lookup"><span data-stu-id="19f72-258">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="19f72-259">Vérifiez qu’aucune règle de refus d’accès n’est configurée pour l’identité de l’utilisateur du pool d’applications sur les répertoires *C:\Program Files\dotnet* et d’application.</span><span class="sxs-lookup"><span data-stu-id="19f72-259">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="19f72-260">Peut-être qu’un déploiement dépendant du framework a été déployé et que .NET Core a été installé sans redémarrage d’IIS.</span><span class="sxs-lookup"><span data-stu-id="19f72-260">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="19f72-261">Redémarrez le serveur ou IIS en exécutant **net stop was /y** suivi de **net start w3svc** à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="19f72-261">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="19f72-262">Peut-être qu’un déploiement dépendant du framework a été déployé sans que le runtime .NET Core soit installé sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="19f72-262">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="19f72-263">Si le runtime .NET Core n’a pas été installé, exécutez le **programme d’installation du bundle d’hébergement .NET Core** sur le système.</span><span class="sxs-lookup"><span data-stu-id="19f72-263">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="19f72-264">Programme d’installation du bundle d’hébergement .NET Core actuel (téléchargement direct)</span><span class="sxs-lookup"><span data-stu-id="19f72-264">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="19f72-265">Pour plus d’informations, consultez [Installer le bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="19f72-265">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="19f72-266">Si un runtime spécifique est nécessaire, téléchargez-le à partir des [archives de téléchargement .NET](https://dotnet.microsoft.com/download/archives), puis installez-le sur le système.</span><span class="sxs-lookup"><span data-stu-id="19f72-266">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="19f72-267">Terminez l’installation en redémarrant le système ou IIS en exécutant **net stop was /y** suivi de **net start w3svc** à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="19f72-267">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="19f72-268">Un déploiement dépendant du framework a peut-être été déployé et *Microsoft Visual C++ 2015 Redistributable (x64)* n’est pas installé sur le système.</span><span class="sxs-lookup"><span data-stu-id="19f72-268">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="19f72-269">Obtenez un programme d’installation à partir du [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="19f72-269">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="19f72-270">Arguments incorrects de l’élément \<aspNetCore></span><span class="sxs-lookup"><span data-stu-id="19f72-270">Incorrect arguments of \<aspNetCore> element</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="19f72-271">**Navigateur :** Erreur HTTP 500.0 - Échec du chargement du gestionnaire in-process d’ANCM</span><span class="sxs-lookup"><span data-stu-id="19f72-271">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="19f72-272">**Journal des applications :** Échec de l’appel de hostfxr pour la recherche du gestionnaire de requêtes in-process. Dépendances natives introuvables.</span><span class="sxs-lookup"><span data-stu-id="19f72-272">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="19f72-273">Cela signifie très probablement que l’application est mal configurée. Vérifiez les versions de Microsoft.NetCore.App et Microsoft.AspNetCore.App ciblées par l’application et installées sur la machine.</span><span class="sxs-lookup"><span data-stu-id="19f72-273">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="19f72-274">Le gestionnaire de requêtes in-process est introuvable.</span><span class="sxs-lookup"><span data-stu-id="19f72-274">Could not find inprocess request handler.</span></span> <span data-ttu-id="19f72-275">Sortie capturée à partir de l’appel de hostfxr : Aviez-vous l’intention d’exécuter les commandes du kit SDK dotnet ?</span><span class="sxs-lookup"><span data-stu-id="19f72-275">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="19f72-276">Installez le kit SDK dotnet à partir de : https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Échec du démarrage de l’application « /LM/W3SVC/3/ROOT ». Code d’erreur : « 0x8000ffff ».</span><span class="sxs-lookup"><span data-stu-id="19f72-276">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="19f72-277">**Journal stdout du module ASP.NET Core :** Aviez-vous l’intention d’exécuter les commandes du kit SDK dotnet ?</span><span class="sxs-lookup"><span data-stu-id="19f72-277">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="19f72-278">Installez le kit SDK dotnet à partir de : https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="19f72-278">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="19f72-279">**Journal de débogage du module ASP.NET Core :** Échec de l’appel de hostfxr pour la recherche du gestionnaire de requêtes in-process. Dépendances natives introuvables.</span><span class="sxs-lookup"><span data-stu-id="19f72-279">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="19f72-280">Cela signifie très probablement que l’application est mal configurée. Vérifiez les versions de Microsoft.NetCore.App et Microsoft.AspNetCore.App ciblées par l’application et installées sur la machine.</span><span class="sxs-lookup"><span data-stu-id="19f72-280">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="19f72-281">HRESULT incorrect retourné : 0x8000ffff Le gestionnaire de requêtes in-process est introuvable.</span><span class="sxs-lookup"><span data-stu-id="19f72-281">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="19f72-282">Sortie capturée à partir de l’appel de hostfxr : Aviez-vous l’intention d’exécuter les commandes du kit SDK dotnet ?</span><span class="sxs-lookup"><span data-stu-id="19f72-282">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="19f72-283">Installez le kit SDK dotnet à partir de : https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 HRESULT incorrect retourné : 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="19f72-283">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="19f72-284">**Navigateur :** Erreur HTTP 502.5 - Échec du processus</span><span class="sxs-lookup"><span data-stu-id="19f72-284">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="19f72-285">**Journal des applications :** L’application « MACHINE/WEBROOT/APPHOST/{ASSEMBLY} » ayant pour racine physique « C:\{CHEMIN}\' » n’a pas pu démarrer le processus à l’aide de la ligne de commande « dotnet .\{ASSEMBLY}.dll ». Code d’erreur = 0x80004005 : 80008081.</span><span class="sxs-lookup"><span data-stu-id="19f72-285">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="19f72-286">**Journal stdout du module ASP.NET Core :** L’application à exécuter n’existe pas : « CHEMIN\{ASSEMBLY}.dll »</span><span class="sxs-lookup"><span data-stu-id="19f72-286">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

::: moniker-end

<span data-ttu-id="19f72-287">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="19f72-287">Troubleshooting:</span></span>

* <span data-ttu-id="19f72-288">Vérifiez que l’application s’exécute localement sur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="19f72-288">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="19f72-289">Un échec de processus peut être dû à un problème au niveau de l’application.</span><span class="sxs-lookup"><span data-stu-id="19f72-289">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="19f72-290">Pour plus d'informations, consultez <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="19f72-290">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="19f72-291">Examinez l’attribut *arguments* de l’élément `<aspNetCore>` dans *web.config* afin de vérifier (a) qu’il s’agit de `.\{ASSEMBLY}.dll` pour un déploiement dépendant du framework, ou (b) qu’il est absent ou qu’il s’agit d’une chaîne vide (`arguments=""`) ou d’une liste d’arguments de l’application (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) pour un déploiement autonome.</span><span class="sxs-lookup"><span data-stu-id="19f72-291">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="19f72-292">Framework partagé .NET Core manquant</span><span class="sxs-lookup"><span data-stu-id="19f72-292">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="19f72-293">**Navigateur :** Erreur HTTP 500.0 - Échec du chargement du gestionnaire in-process d’ANCM</span><span class="sxs-lookup"><span data-stu-id="19f72-293">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="19f72-294">**Journal des applications :** Échec de l’appel de hostfxr pour la recherche du gestionnaire de requêtes in-process. Dépendances natives introuvables.</span><span class="sxs-lookup"><span data-stu-id="19f72-294">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="19f72-295">Cela signifie très probablement que l’application est mal configurée. Vérifiez les versions de Microsoft.NetCore.App et Microsoft.AspNetCore.App ciblées par l’application et installées sur la machine.</span><span class="sxs-lookup"><span data-stu-id="19f72-295">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="19f72-296">Le gestionnaire de requêtes in-process est introuvable.</span><span class="sxs-lookup"><span data-stu-id="19f72-296">Could not find inprocess request handler.</span></span> <span data-ttu-id="19f72-297">Sortie capturée à partir de l’appel de hostfxr : Il n’a pas été possible de localiser une version compatible du framework.</span><span class="sxs-lookup"><span data-stu-id="19f72-297">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="19f72-298">Le framework spécifié « Microsoft.AspNetCore.App », version « {VERSION} » est introuvable.</span><span class="sxs-lookup"><span data-stu-id="19f72-298">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="19f72-299">Échec du démarrage de l’application « /LM/W3SVC/5/ROOT ». Code d’erreur : 0x8000ffff.</span><span class="sxs-lookup"><span data-stu-id="19f72-299">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="19f72-300">**Journal stdout du module ASP.NET Core :** Il n’a pas été possible de localiser une version compatible du framework.</span><span class="sxs-lookup"><span data-stu-id="19f72-300">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="19f72-301">Le framework spécifié « Microsoft.AspNetCore.App », version « {VERSION} » est introuvable.</span><span class="sxs-lookup"><span data-stu-id="19f72-301">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="19f72-302">**Journal de débogage du module ASP.NET Core :** HRESULT incorrect retourné : 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="19f72-302">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

<span data-ttu-id="19f72-303">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="19f72-303">Troubleshooting:</span></span>

<span data-ttu-id="19f72-304">Pour un déploiement dépendant du framework, vérifiez que le runtime approprié est installé sur le système.</span><span class="sxs-lookup"><span data-stu-id="19f72-304">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="19f72-305">Pool d’applications arrêté</span><span class="sxs-lookup"><span data-stu-id="19f72-305">Stopped Application Pool</span></span>

* <span data-ttu-id="19f72-306">**Navigateur :** 503 Service indisponible</span><span class="sxs-lookup"><span data-stu-id="19f72-306">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="19f72-307">**Journal des applications :** Entrée interdite</span><span class="sxs-lookup"><span data-stu-id="19f72-307">**Application Log:** No entry</span></span>

* <span data-ttu-id="19f72-308">**Journal stdout du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="19f72-308">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="19f72-309">**Journal de débogage du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="19f72-309">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="19f72-310">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="19f72-310">Troubleshooting:</span></span>

<span data-ttu-id="19f72-311">Vérifiez que le pool d’applications n’est pas à l’état *Arrêté*.</span><span class="sxs-lookup"><span data-stu-id="19f72-311">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="19f72-312">La sous-application inclut une section \<handlers></span><span class="sxs-lookup"><span data-stu-id="19f72-312">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="19f72-313">**Navigateur :** HTTP Erreur 500.19 - Erreur de serveur interne</span><span class="sxs-lookup"><span data-stu-id="19f72-313">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="19f72-314">**Journal des applications :** Entrée interdite</span><span class="sxs-lookup"><span data-stu-id="19f72-314">**Application Log:** No entry</span></span>

* <span data-ttu-id="19f72-315">**Journal stdout du module ASP.NET Core :** Le fichier journal de l’application racine est créé et indique un fonctionnement normal.</span><span class="sxs-lookup"><span data-stu-id="19f72-315">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="19f72-316">Le fichier journal de la sous-application n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="19f72-316">The sub-app's log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="19f72-317">**Journal de débogage du module ASP.NET Core :** Le fichier journal de l’application racine est créé et indique un fonctionnement normal.</span><span class="sxs-lookup"><span data-stu-id="19f72-317">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="19f72-318">Le fichier journal de la sous-application n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="19f72-318">The sub-app's log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="19f72-319">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="19f72-319">Troubleshooting:</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="19f72-320">Vérifiez que le fichier *web.config* de la sous-application n’inclut pas de section `<handlers>` ou que la sous-application n’hérite pas des gestionnaires de l’application parente.</span><span class="sxs-lookup"><span data-stu-id="19f72-320">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="19f72-321">La section `<system.webServer>` de l’application parente de *web.config* est placée à l’intérieur d’un élément `<location>`.</span><span class="sxs-lookup"><span data-stu-id="19f72-321">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="19f72-322">La propriété <xref:System.Configuration.SectionInformation.InheritInChildApplications*> a la valeur `false` pour indiquer que les paramètres spécifiés dans l’élément [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) ne sont pas hérités par les applications situées dans un sous-répertoire de l’application parente.</span><span class="sxs-lookup"><span data-stu-id="19f72-322">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="19f72-323">Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="19f72-323">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="19f72-324">Vérifiez que le fichier *web.config* de la sous-application n’inclut pas de section `<handlers>`.</span><span class="sxs-lookup"><span data-stu-id="19f72-324">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

::: moniker-end

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="19f72-325">Chemin du journal stdout incorrect</span><span class="sxs-lookup"><span data-stu-id="19f72-325">stdout log path incorrect</span></span>

* <span data-ttu-id="19f72-326">**Navigateur :** L’application répond normalement.</span><span class="sxs-lookup"><span data-stu-id="19f72-326">**Browser:** The app responds normally.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="19f72-327">**Journal des applications :** Impossible de démarrer la redirection de stdout dans C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="19f72-327">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="19f72-328">Message d'exception : HRESULT 0x80070005 retourné sur {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span><span class="sxs-lookup"><span data-stu-id="19f72-328">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="19f72-329">Impossible d’arrêter la redirection de stdout dans C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="19f72-329">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="19f72-330">Message d'exception : HRESULT 0x80070002 retourné sur {CHEMIN}.</span><span class="sxs-lookup"><span data-stu-id="19f72-330">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="19f72-331">Impossible de démarrer la redirection de stdout dans {CHEMIN}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="19f72-331">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="19f72-332">**Journal stdout du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="19f72-332">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="19f72-333">**Journal de débogage du module ASP.NET Core :** Impossible de démarrer la redirection de stdout dans C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="19f72-333">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="19f72-334">Message d'exception : HRESULT 0x80070005 retourné sur {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span><span class="sxs-lookup"><span data-stu-id="19f72-334">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="19f72-335">Impossible d’arrêter la redirection de stdout dans C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="19f72-335">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="19f72-336">Message d'exception : HRESULT 0x80070002 retourné sur {CHEMIN}.</span><span class="sxs-lookup"><span data-stu-id="19f72-336">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="19f72-337">Impossible de démarrer la redirection de stdout dans {CHEMIN}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="19f72-337">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="19f72-338">**Journal des applications :** Avertissement : Impossible de créer stdoutLogFile \\?\{CHEMIN}\path_doesnt_exist\stdout_{ID_DE_PROCESSUS}_{HORODATAGE}.log. Code d’erreur = -2147024893.</span><span class="sxs-lookup"><span data-stu-id="19f72-338">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="19f72-339">**Journal stdout du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="19f72-339">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="19f72-340">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="19f72-340">Troubleshooting:</span></span>

* <span data-ttu-id="19f72-341">Le chemin `stdoutLogFile` spécifié dans l’élément `<aspNetCore>` de *web.config* n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="19f72-341">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="19f72-342">Pour plus d’informations, consultez [Module ASP.NET Core : Création et redirection de journal](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span><span class="sxs-lookup"><span data-stu-id="19f72-342">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="19f72-343">L’utilisateur du pool d’applications ne dispose pas d’un accès en écriture sur le chemin du journal stdout.</span><span class="sxs-lookup"><span data-stu-id="19f72-343">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="19f72-344">Problème général lié à la configuration d’application</span><span class="sxs-lookup"><span data-stu-id="19f72-344">Application configuration general issue</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="19f72-345">**Navigateur :** Erreur HTTP 500.0 - Échec du chargement du gestionnaire in-process d’ANCM **--OU--** Erreur HTTP 500.30 - Échec du démarrage in-process d’ANCM</span><span class="sxs-lookup"><span data-stu-id="19f72-345">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="19f72-346">**Journal des applications :** Variable</span><span class="sxs-lookup"><span data-stu-id="19f72-346">**Application Log:** Variable</span></span>

* <span data-ttu-id="19f72-347">**Journal stdout du module ASP.NET Core :** Le fichier journal est créé mais il est vide ou a été créé avec des entrées normales jusqu’au point d’échec de l’application.</span><span class="sxs-lookup"><span data-stu-id="19f72-347">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="19f72-348">**Journal de débogage du module ASP.NET Core :** Variable</span><span class="sxs-lookup"><span data-stu-id="19f72-348">**ASP.NET Core Module Debug Log:** Variable</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="19f72-349">**Navigateur :** Erreur HTTP 502.5 - Échec du processus</span><span class="sxs-lookup"><span data-stu-id="19f72-349">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="19f72-350">**Journal des applications :** L’application « MACHINE/WEBROOT/APPHOST/{ASSEMBLY} » ayant pour racine physique « C:\{CHEMIN}\' » a créé un processus à l’aide de la ligne de commande « C:\{CHEMIN}\{ASSEMBLY}.{exe|dll} ». Toutefois, l’application a planté, n’a pas répondu ou n’a pas écouté sur le port indiqué « {PORT} ». Code d’erreur = « {CODE D’ERREUR} »</span><span class="sxs-lookup"><span data-stu-id="19f72-350">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="19f72-351">**Journal stdout du module ASP.NET Core :** Le fichier journal est créé mais il est vide.</span><span class="sxs-lookup"><span data-stu-id="19f72-351">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="19f72-352">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="19f72-352">Troubleshooting:</span></span>

<span data-ttu-id="19f72-353">Le processus n’a pas pu démarrer, probablement en raison d’un problème de configuration ou de programmation d’application.</span><span class="sxs-lookup"><span data-stu-id="19f72-353">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="19f72-354">Pour plus d’informations, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="19f72-354">For more information, see the following topics:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:test/troubleshoot>
