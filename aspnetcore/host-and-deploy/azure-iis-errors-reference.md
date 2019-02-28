---
title: Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core
author: guardrex
description: Obtenez des conseils de résolution de problèmes pour les erreurs courantes liées à l’hébergement d’applications ASP.NET Core sur Azure Apps Service et IIS.
ms.author: riande
ms.custom: mvc
ms.date: 02/21/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: d1cdac4d27ee1bc3ebb4329c1bbd3bdacb34a58c
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56743945"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="bfa8f-103">Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bfa8f-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="bfa8f-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bfa8f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bfa8f-105">Cette rubrique offre des conseils de résolution de problèmes pour les erreurs courantes liées à l’hébergement d’applications ASP.NET Core sur Azure Apps Service et IIS.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-105">This topic offers troubleshooting advice for common errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="bfa8f-106">Collectez les informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-106">Collect the following information:</span></span>

* <span data-ttu-id="bfa8f-107">Comportement du navigateur (code d’état et message d’erreur)</span><span class="sxs-lookup"><span data-stu-id="bfa8f-107">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="bfa8f-108">Entrées du journal des événements de l’application</span><span class="sxs-lookup"><span data-stu-id="bfa8f-108">Application Event Log entries</span></span>
  * <span data-ttu-id="bfa8f-109">Azure App Service &ndash; Consultez <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-109">Azure App Service &ndash; See <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
  * <span data-ttu-id="bfa8f-110">IIS</span><span class="sxs-lookup"><span data-stu-id="bfa8f-110">IIS</span></span>
    1. <span data-ttu-id="bfa8f-111">Sélectionnez **Démarrer** dans le menu **Windows**, tapez *Observateur d’événements*, puis appuyez sur **Entrée**.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-111">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="bfa8f-112">Une fois l’**Observateur d’événements** ouvert, développez **Journaux Windows** > **Application** dans la barre latérale.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-112">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="bfa8f-113">Entrées de journal stdout et de débogage du module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bfa8f-113">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="bfa8f-114">Azure App Service &ndash; Consultez <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-114">Azure App Service &ndash; See <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
  * <span data-ttu-id="bfa8f-115">IIS &ndash; Suivez les instructions données dans les sections [Création et redirection de journal](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) et [Journaux de diagnostic améliorés](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) de la rubrique Module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-115">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="bfa8f-116">Comparez les informations d’erreur aux erreurs courantes suivantes.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-116">Compare error information to the following common errors.</span></span> <span data-ttu-id="bfa8f-117">Si vous trouvez une correspondance, suivez les conseils de dépannage.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-117">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="bfa8f-118">La liste d’erreurs de cette rubrique n’est pas exhaustive.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-118">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="bfa8f-119">Si vous rencontrez une erreur non listée ici, ouvrez un nouveau problème à l’aide du bouton de **Commentaires sur le contenu** situé au bas de cette rubrique, puis fournissez des instructions détaillées sur la manière de reproduire l’erreur.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-119">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="bfa8f-120">Le programme d’installation ne parvient pas à obtenir VC++ Redistributable</span><span class="sxs-lookup"><span data-stu-id="bfa8f-120">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="bfa8f-121">**Exception du programme d’installation :** 0x80072efd **--OU--** 0x80072f76 - Erreur non spécifiée</span><span class="sxs-lookup"><span data-stu-id="bfa8f-121">**Installer Exception:** 0x80072efd **--OR--** 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="bfa8f-122">**Exception du journal d’installation&#8224; :** Erreur 0x80072efd **--OU--** 0x80072f76 : Échec de l’exécution du package EXE</span><span class="sxs-lookup"><span data-stu-id="bfa8f-122">**Installer Log Exception&#8224;:** Error 0x80072efd **--OR--** 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="bfa8f-123">&#8224;Le journal se trouve sur *C:\Users\{UTILISATEUR}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{HORODATAGE}.log*.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-123">&#8224;The log is located at *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span></span>

<span data-ttu-id="bfa8f-124">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-124">Troubleshooting:</span></span>

<span data-ttu-id="bfa8f-125">Si le système n’a pas accès à Internet au moment de l’[installation du bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), cette exception se produit quand le programme d’installation ne peut pas obtenir *Microsoft Visual C++ 2015 Redistributable*.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-125">If the system doesn't have Internet access while [installing the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="bfa8f-126">Obtenez un programme d’installation à partir du [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="bfa8f-126">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="bfa8f-127">En cas d’échec du programme d’installation, le serveur risque de ne pas recevoir le runtime .NET Core nécessaire à l’hébergement d’un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="bfa8f-127">If the installer fails, the server may not receive the .NET Core runtime required to host a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span> <span data-ttu-id="bfa8f-128">En cas d’hébergement d’un déploiement dépendant du framework, vérifiez que le runtime est installé dans **Programmes et fonctionnalités** ou **Applications et fonctionnalités**.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-128">If hosting an FDD, confirm that the runtime is installed in **Programs & Features** or **Apps & features**.</span></span> <span data-ttu-id="bfa8f-129">Si un runtime spécifique est nécessaire, téléchargez-le à partir des [archives de téléchargement .NET](https://dotnet.microsoft.com/download/archives), puis installez-le sur le système.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-129">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="bfa8f-130">Après avoir installé le runtime, redémarrez le système ou IIS en exécutant **net stop was /y** suivi de **net start w3svc** à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-130">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="bfa8f-131">La mise à niveau du système d’exploitation a supprimé le Module ASP.NET Core 32 bits</span><span class="sxs-lookup"><span data-stu-id="bfa8f-131">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="bfa8f-132">**Journal des applications :** Le fichier DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** du module n’a pas pu se charger.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-132">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="bfa8f-133">Les données sont erronées.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-133">The data is the error.</span></span>

<span data-ttu-id="bfa8f-134">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-134">Troubleshooting:</span></span>

<span data-ttu-id="bfa8f-135">Les fichiers autres que les fichiers de système d’exploitation dans le répertoire **C:\Windows\SysWOW64\inetsrv** ne sont pas conservés pendant la mise à niveau du système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-135">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="bfa8f-136">Si le module ASP.NET Core est installé avant la mise à niveau d’un système d’exploitation et si un pool d’applications est exécuté en mode 32 bits après la mise à niveau du système d’exploitation, ce problème se produit.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-136">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="bfa8f-137">Après une mise à niveau du système d’exploitation, réparez le Module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-137">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="bfa8f-138">Consultez [Installer le bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="bfa8f-138">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="bfa8f-139">Sélectionnez **Réparer** quand le programme d’installation est exécuté.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-139">Select **Repair** when the installer is run.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="bfa8f-140">Une application x86 est déployée mais le pool d’applications n’est pas activé pour les applications 32 bits</span><span class="sxs-lookup"><span data-stu-id="bfa8f-140">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="bfa8f-141">**Navigateur :** Erreur HTTP 500.30 - Échec du démarrage in-process d’ANCM</span><span class="sxs-lookup"><span data-stu-id="bfa8f-141">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="bfa8f-142">**Journal des applications :** L’application « /LM/W3SVC/5/ROOT » ayant pour racine physique « {PATH} » a rencontré une exception managée inattendue. Code d’exception = « 0xe0434352 ».</span><span class="sxs-lookup"><span data-stu-id="bfa8f-142">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="bfa8f-143">Pour plus d’informations, consultez les journaux stderr.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-143">Please check the stderr logs for more information.</span></span> <span data-ttu-id="bfa8f-144">L’application « /LM/W3SVC/5/ROOT » ayant pour racine physique « {PATH} » n’a pas pu charger clr et l’application managée.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-144">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="bfa8f-145">Sortie prématurée du thread de travail CLR</span><span class="sxs-lookup"><span data-stu-id="bfa8f-145">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="bfa8f-146">**Journal stdout du module ASP.NET Core :** Le fichier journal est créé mais il est vide.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-146">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bfa8f-147">**Journal de débogage du module ASP.NET Core :** HRESULT incorrect retourné : 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="bfa8f-147">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

<span data-ttu-id="bfa8f-148">Ce scénario est intercepté par le kit SDK au moment de la publication d’une application autonome.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-148">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="bfa8f-149">Le kit SDK génère une erreur si le RID ne correspond pas à la cible de la plateforme (par exemple, un RID `win10-x64` avec `<PlatformTarget>x86</PlatformTarget>` dans le fichier projet).</span><span class="sxs-lookup"><span data-stu-id="bfa8f-149">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="bfa8f-150">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-150">Troubleshooting:</span></span>

<span data-ttu-id="bfa8f-151">Pour un déploiement dépendant du framework x86 (`<PlatformTarget>x86</PlatformTarget>`), activez le pool d’applications IIS pour les applications 32 bits.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-151">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="bfa8f-152">Dans le Gestionnaire IIS, ouvrez les **Paramètres avancés** du pool d’applications, puis affectez à l’option **Activer les applications 32 bits** la valeur **Vrai**.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-152">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="bfa8f-153">Conflits de plateforme avec RID</span><span class="sxs-lookup"><span data-stu-id="bfa8f-153">Platform conflicts with RID</span></span>

* <span data-ttu-id="bfa8f-154">**Navigateur :** Erreur HTTP 502.5 - Échec du processus</span><span class="sxs-lookup"><span data-stu-id="bfa8f-154">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bfa8f-155">**Journal des applications :** L’application « MACHINE/WEBROOT/APPHOST/{ASSEMBLY} » ayant pour racine physique « C:\{CHEMIN}\' » n’a pas pu démarrer le processus à l’aide de la ligne de commande « C:\{CHEMIN}{ASSEMBLY}.{exe|dll} ». Code d’erreur = « 0x80004005 : ff ».</span><span class="sxs-lookup"><span data-stu-id="bfa8f-155">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="bfa8f-156">**Journal stdout du module ASP.NET Core :** Exception non prise en charge : System.BadImageFormatException : Impossible de charger le fichier ou l’assembly « {ASSEMBLY}.dll ».</span><span class="sxs-lookup"><span data-stu-id="bfa8f-156">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="bfa8f-157">Tentative de chargement d’un programme au format incorrect.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-157">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="bfa8f-158">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-158">Troubleshooting:</span></span>

* <span data-ttu-id="bfa8f-159">Vérifiez que l’application s’exécute localement sur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-159">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="bfa8f-160">Un échec de processus peut être dû à un problème au niveau de l’application.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-160">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="bfa8f-161">Pour plus d’informations, consultez [Résoudre les problèmes (IIS)](xref:host-and-deploy/iis/troubleshoot) ou [Résoudre les problèmes (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="bfa8f-161">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="bfa8f-162">Si cette exception se produit pour un déploiement d’applications Azure pendant la mise à niveau d’une application et le déploiement de nouveaux assemblys, supprimez manuellement tous les fichiers du déploiement précédent.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-162">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="bfa8f-163">Le fait de laisser des assemblys incompatibles peut provoquer une exception `System.BadImageFormatException` lors du déploiement d’une application mise à niveau.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-163">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="bfa8f-164">Point de terminaison d’URI incorrect ou site web arrêté</span><span class="sxs-lookup"><span data-stu-id="bfa8f-164">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="bfa8f-165">**Navigateur :** ERR_CONNECTION_REFUSED **--OU--** Connexion impossible</span><span class="sxs-lookup"><span data-stu-id="bfa8f-165">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="bfa8f-166">**Journal des applications :** Entrée interdite</span><span class="sxs-lookup"><span data-stu-id="bfa8f-166">**Application Log:** No entry</span></span>

* <span data-ttu-id="bfa8f-167">**Journal stdout du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-167">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bfa8f-168">**Journal de débogage du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-168">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="bfa8f-169">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-169">Troubleshooting:</span></span>

* <span data-ttu-id="bfa8f-170">Vérifiez que le point de terminaison d’URI approprié de l’application est en cours d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-170">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="bfa8f-171">Vérifiez les liaisons.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-171">Check the bindings.</span></span>

* <span data-ttu-id="bfa8f-172">Vérifiez que le site web IIS n’est pas à l’état *Arrêté*.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-172">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="bfa8f-173">Fonctionnalités de serveur W3SVC ou CoreWebEngine désactivées</span><span class="sxs-lookup"><span data-stu-id="bfa8f-173">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="bfa8f-174">**Exception de système d’exploitation :** Les fonctionnalités CoreWebEngine et W3SVC d’IIS 7.0 doivent être installées pour permettre l’utilisation du module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-174">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="bfa8f-175">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-175">Troubleshooting:</span></span>

<span data-ttu-id="bfa8f-176">Vérifiez que le rôle et les fonctionnalités appropriés sont activés.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-176">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="bfa8f-177">Consultez [Configuration d’IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="bfa8f-177">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="bfa8f-178">Chemin physique de site web incorrect ou application manquante</span><span class="sxs-lookup"><span data-stu-id="bfa8f-178">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="bfa8f-179">**Navigateur :** 403 Interdit - Accès refusé **--OU--** 403.14 Interdit - Le serveur web est configuré pour ne pas afficher le contenu de ce répertoire.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-179">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="bfa8f-180">**Journal des applications :** Entrée interdite</span><span class="sxs-lookup"><span data-stu-id="bfa8f-180">**Application Log:** No entry</span></span>

* <span data-ttu-id="bfa8f-181">**Journal stdout du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-181">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bfa8f-182">**Journal de débogage du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-182">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="bfa8f-183">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-183">Troubleshooting:</span></span>

<span data-ttu-id="bfa8f-184">Consultez les **Paramètres de base** du site web IIS et le dossier d’application physique.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-184">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="bfa8f-185">Vérifiez que l’application est dans le dossier sur le **chemin physique** du site web IIS.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-185">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="bfa8f-186">Rôle incorrect, module ASP.NET Core non installé ou autorisations incorrectes</span><span class="sxs-lookup"><span data-stu-id="bfa8f-186">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="bfa8f-187">**Navigateur :** 500.19 Erreur de serveur interne - Impossible d’accéder à la page demandée, car les données de configuration associées à la page sont non valides.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-187">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="bfa8f-188">**--OU--** Impossible d’afficher cette page</span><span class="sxs-lookup"><span data-stu-id="bfa8f-188">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="bfa8f-189">**Journal des applications :** Entrée interdite</span><span class="sxs-lookup"><span data-stu-id="bfa8f-189">**Application Log:** No entry</span></span>

* <span data-ttu-id="bfa8f-190">**Journal stdout du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-190">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bfa8f-191">**Journal de débogage du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-191">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="bfa8f-192">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-192">Troubleshooting:</span></span>

* <span data-ttu-id="bfa8f-193">Vérifiez que le rôle approprié est activé.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-193">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="bfa8f-194">Consultez [Configuration d’IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="bfa8f-194">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="bfa8f-195">Ouvrez **Programmes et fonctionnalités** ou **Applications et fonctionnalités**, puis vérifiez que **Windows Server Hosting** est installé.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-195">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="bfa8f-196">Si **Windows Server Hosting** ne figure pas dans la liste des programmes installés, téléchargez et installez le bundle d’hébergement .NET Core.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-196">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="bfa8f-197">Programme d’installation du bundle d’hébergement .NET Core actuel (téléchargement direct)</span><span class="sxs-lookup"><span data-stu-id="bfa8f-197">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="bfa8f-198">Pour plus d’informations, consultez [Installer le bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="bfa8f-198">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="bfa8f-199">Vérifiez que **Pool d’applications** > **Modèle de processus** > **Identité** a la valeur **ApplicationPoolIdentity** ou que l’identité personnalisée dispose des autorisations appropriées pour accéder au dossier de déploiement de l’application.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-199">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="bfa8f-200">Si vous avez désinstallé le bundle d’hébergement ASP.NET Core et installé une version antérieure du bundle d’hébergement, le fichier *applicationHost.config* ne contient pas de section pour le module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-200">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="bfa8f-201">Ouvrez *applicationHost.config* sur *%windir%/System32/inetsrv/config* et recherchez le groupe de sections `<configuration><configSections><sectionGroup name="system.webServer">`.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-201">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="bfa8f-202">Si la section pour le module ASP.NET Core ne se trouve pas dans le groupe de sections, ajoutez l’élément de section :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-202">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```
  
  <span data-ttu-id="bfa8f-203">Sinon, installez la dernière version du bundle d’hébergement ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-203">Alternatively, install the lastest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="bfa8f-204">La dernière version est rétrocompatible avec les applications ASP.NET Core prises en charge.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-204">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="bfa8f-205">processPath incorrect, variable de chemin manquante, bundle d’hébergement non installé, système/IIS non redémarré, VC++ Redistributable non installé ou violation d’accès dotnet.exe</span><span class="sxs-lookup"><span data-stu-id="bfa8f-205">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bfa8f-206">**Navigateur :** Erreur HTTP 500.0 - Échec du chargement du gestionnaire in-process d’ANCM</span><span class="sxs-lookup"><span data-stu-id="bfa8f-206">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="bfa8f-207">**Journal des applications :** L’application « MACHINE/WEBROOT/APPHOST/{ASSEMBLY} » ayant pour racine physique « C:\{CHEMIN}\' » n’a pas pu démarrer le processus à l’aide de la ligne de commande « {...} »</span><span class="sxs-lookup"><span data-stu-id="bfa8f-207">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="bfa8f-208">Code d’erreur = 0x80070002 : 0.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-208">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="bfa8f-209">L’application « {PATH} » n’a pas pu démarrer.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-209">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="bfa8f-210">L’exécutable est introuvable sur « {PATH} ».</span><span class="sxs-lookup"><span data-stu-id="bfa8f-210">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="bfa8f-211">Échec du démarrage de l’application « /LM/W3SVC/2/ROOT ». Code d’erreur : 0x8007023e.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-211">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="bfa8f-212">**Journal stdout du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-212">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="bfa8f-213">**Journal de débogage du module ASP.NET Core :** Journal des événements : L’application « {PATH} » n’a pas pu démarrer.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-213">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="bfa8f-214">L’exécutable est introuvable sur « {PATH} ».</span><span class="sxs-lookup"><span data-stu-id="bfa8f-214">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="bfa8f-215">HRESULT incorrect retourné : 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="bfa8f-215">Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="bfa8f-216">**Navigateur :** Erreur HTTP 502.5 - Échec du processus</span><span class="sxs-lookup"><span data-stu-id="bfa8f-216">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bfa8f-217">**Journal des applications :** L’application « MACHINE/WEBROOT/APPHOST/{ASSEMBLY} » ayant pour racine physique « C:\{CHEMIN}\' » n’a pas pu démarrer le processus à l’aide de la ligne de commande « {...} »</span><span class="sxs-lookup"><span data-stu-id="bfa8f-217">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="bfa8f-218">Code d’erreur = 0x80070002 : 0.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-218">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="bfa8f-219">**Journal stdout du module ASP.NET Core :** Le fichier journal est créé mais il est vide.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-219">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="bfa8f-220">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-220">Troubleshooting:</span></span>

* <span data-ttu-id="bfa8f-221">Vérifiez que l’application s’exécute localement sur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-221">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="bfa8f-222">Un échec de processus peut être dû à un problème au niveau de l’application.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-222">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="bfa8f-223">Pour plus d’informations, consultez [Résoudre les problèmes (IIS)](xref:host-and-deploy/iis/troubleshoot) ou [Résoudre les problèmes (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="bfa8f-223">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="bfa8f-224">Examinez l’attribut *processPath* de l’élément `<aspNetCore>` dans *web.config* afin de vérifier qu’il s’agit de `dotnet` pour un déploiement dépendant du framework ou de `.\{ASSEMBLY}.exe` pour un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="bfa8f-224">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="bfa8f-225">Pour un déploiement dépendant du framework, *dotnet.exe* peut ne pas être accessible via les paramètres PATH.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-225">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="bfa8f-226">Vérifiez que *C:\Program Files\dotnet\\* existe dans les paramètres PATH du système.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-226">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="bfa8f-227">Dans le cas d’un déploiement dépendant du framework, *dotnet.exe* risque de ne pas être accessible pour l’identité de l’utilisateur du pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-227">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="bfa8f-228">Vérifiez que l’identité de l’utilisateur du pool d’applications a accès au répertoire *C:\Program Files\dotnet*.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-228">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="bfa8f-229">Vérifiez qu’aucune règle de refus d’accès n’est configurée pour l’identité de l’utilisateur du pool d’applications sur les répertoires *C:\Program Files\dotnet* et d’application.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-229">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="bfa8f-230">Peut-être qu’un déploiement dépendant du framework a été déployé et que .NET Core a été installé sans redémarrage d’IIS.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-230">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="bfa8f-231">Redémarrez le serveur ou IIS en exécutant **net stop was /y** suivi de **net start w3svc** à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-231">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="bfa8f-232">Peut-être qu’un déploiement dépendant du framework a été déployé sans que le runtime .NET Core soit installé sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-232">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="bfa8f-233">Si le runtime .NET Core n’a pas été installé, exécutez le **programme d’installation du bundle d’hébergement .NET Core** sur le système.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-233">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="bfa8f-234">Programme d’installation du bundle d’hébergement .NET Core actuel (téléchargement direct)</span><span class="sxs-lookup"><span data-stu-id="bfa8f-234">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="bfa8f-235">Pour plus d’informations, consultez [Installer le bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="bfa8f-235">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="bfa8f-236">Si un runtime spécifique est nécessaire, téléchargez-le à partir des [archives de téléchargement .NET](https://dotnet.microsoft.com/download/archives), puis installez-le sur le système.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-236">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="bfa8f-237">Terminez l’installation en redémarrant le système ou IIS en exécutant **net stop was /y** suivi de **net start w3svc** à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-237">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="bfa8f-238">Un déploiement dépendant du framework a peut-être été déployé et *Microsoft Visual C++ 2015 Redistributable (x64)* n’est pas installé sur le système.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-238">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="bfa8f-239">Obtenez un programme d’installation à partir du [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="bfa8f-239">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="bfa8f-240">Arguments incorrects de l’élément \<aspNetCore></span><span class="sxs-lookup"><span data-stu-id="bfa8f-240">Incorrect arguments of \<aspNetCore> element</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bfa8f-241">**Navigateur :** Erreur HTTP 500.0 - Échec du chargement du gestionnaire in-process d’ANCM</span><span class="sxs-lookup"><span data-stu-id="bfa8f-241">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="bfa8f-242">**Journal des applications :** Échec de l’appel de hostfxr pour la recherche du gestionnaire de requêtes in-process. Dépendances natives introuvables.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-242">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="bfa8f-243">Cela signifie très probablement que l’application est mal configurée. Vérifiez les versions de Microsoft.NetCore.App et Microsoft.AspNetCore.App ciblées par l’application et installées sur la machine.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-243">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="bfa8f-244">Le gestionnaire de requêtes in-process est introuvable.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-244">Could not find inprocess request handler.</span></span> <span data-ttu-id="bfa8f-245">Sortie capturée à partir de l’appel de hostfxr : Aviez-vous l’intention d’exécuter les commandes du kit SDK dotnet ?</span><span class="sxs-lookup"><span data-stu-id="bfa8f-245">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="bfa8f-246">Installez le kit SDK dotnet à partir de : https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Échec du démarrage de l’application « /LM/W3SVC/3/ROOT ». Code d’erreur : « 0x8000ffff ».</span><span class="sxs-lookup"><span data-stu-id="bfa8f-246">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="bfa8f-247">**Journal stdout du module ASP.NET Core :** Aviez-vous l’intention d’exécuter les commandes du kit SDK dotnet ?</span><span class="sxs-lookup"><span data-stu-id="bfa8f-247">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="bfa8f-248">Installez le kit SDK dotnet à partir de : https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="bfa8f-248">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="bfa8f-249">**Journal de débogage du module ASP.NET Core :** Échec de l’appel de hostfxr pour la recherche du gestionnaire de requêtes in-process. Dépendances natives introuvables.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-249">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="bfa8f-250">Cela signifie très probablement que l’application est mal configurée. Vérifiez les versions de Microsoft.NetCore.App et Microsoft.AspNetCore.App ciblées par l’application et installées sur la machine.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-250">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="bfa8f-251">HRESULT incorrect retourné : 0x8000ffff Le gestionnaire de requêtes in-process est introuvable.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-251">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="bfa8f-252">Sortie capturée à partir de l’appel de hostfxr : Aviez-vous l’intention d’exécuter les commandes du kit SDK dotnet ?</span><span class="sxs-lookup"><span data-stu-id="bfa8f-252">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="bfa8f-253">Installez le kit SDK dotnet à partir de : https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 HRESULT incorrect retourné : 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="bfa8f-253">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="bfa8f-254">**Navigateur :** Erreur HTTP 502.5 - Échec du processus</span><span class="sxs-lookup"><span data-stu-id="bfa8f-254">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bfa8f-255">**Journal des applications :** L’application « MACHINE/WEBROOT/APPHOST/{ASSEMBLY} » ayant pour racine physique « C:\{CHEMIN}\' » n’a pas pu démarrer le processus à l’aide de la ligne de commande « dotnet .\{ASSEMBLY}.dll ». Code d’erreur = 0x80004005 : 80008081.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-255">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="bfa8f-256">**Journal stdout du module ASP.NET Core :** L’application à exécuter n’existe pas : « CHEMIN\{ASSEMBLY}.dll »</span><span class="sxs-lookup"><span data-stu-id="bfa8f-256">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

::: moniker-end

<span data-ttu-id="bfa8f-257">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-257">Troubleshooting:</span></span>

* <span data-ttu-id="bfa8f-258">Vérifiez que l’application s’exécute localement sur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-258">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="bfa8f-259">Un échec de processus peut être dû à un problème au niveau de l’application.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-259">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="bfa8f-260">Pour plus d’informations, consultez [Résoudre les problèmes (IIS)](xref:host-and-deploy/iis/troubleshoot) ou [Résoudre les problèmes (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="bfa8f-260">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="bfa8f-261">Examinez l’attribut *arguments* de l’élément `<aspNetCore>` dans *web.config* afin de vérifier (a) qu’il s’agit de `.\{ASSEMBLY}.dll` pour un déploiement dépendant du framework, ou (b) qu’il est absent ou qu’il s’agit d’une chaîne vide (`arguments=""`) ou d’une liste d’arguments de l’application (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) pour un déploiement autonome.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-261">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="bfa8f-262">Framework partagé .NET Core manquant</span><span class="sxs-lookup"><span data-stu-id="bfa8f-262">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="bfa8f-263">**Navigateur :** Erreur HTTP 500.0 - Échec du chargement du gestionnaire in-process d’ANCM</span><span class="sxs-lookup"><span data-stu-id="bfa8f-263">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="bfa8f-264">**Journal des applications :** Échec de l’appel de hostfxr pour la recherche du gestionnaire de requêtes in-process. Dépendances natives introuvables.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-264">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="bfa8f-265">Cela signifie très probablement que l’application est mal configurée. Vérifiez les versions de Microsoft.NetCore.App et Microsoft.AspNetCore.App ciblées par l’application et installées sur la machine.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-265">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="bfa8f-266">Le gestionnaire de requêtes in-process est introuvable.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-266">Could not find inprocess request handler.</span></span> <span data-ttu-id="bfa8f-267">Sortie capturée à partir de l’appel de hostfxr : Il n’a pas été possible de localiser une version compatible du framework.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-267">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="bfa8f-268">Le framework spécifié « Microsoft.AspNetCore.App », version « {VERSION} » est introuvable.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-268">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="bfa8f-269">Échec du démarrage de l’application « /LM/W3SVC/5/ROOT ». Code d’erreur : 0x8000ffff.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-269">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="bfa8f-270">**Journal stdout du module ASP.NET Core :** Il n’a pas été possible de localiser une version compatible du framework.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-270">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="bfa8f-271">Le framework spécifié « Microsoft.AspNetCore.App », version « {VERSION} » est introuvable.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-271">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="bfa8f-272">**Journal de débogage du module ASP.NET Core :** HRESULT incorrect retourné : 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="bfa8f-272">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

<span data-ttu-id="bfa8f-273">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-273">Troubleshooting:</span></span>

<span data-ttu-id="bfa8f-274">Pour un déploiement dépendant du framework, vérifiez que le runtime approprié est installé sur le système.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-274">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="bfa8f-275">Pool d’applications arrêté</span><span class="sxs-lookup"><span data-stu-id="bfa8f-275">Stopped Application Pool</span></span>

* <span data-ttu-id="bfa8f-276">**Navigateur :** 503 Service indisponible</span><span class="sxs-lookup"><span data-stu-id="bfa8f-276">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="bfa8f-277">**Journal des applications :** Entrée interdite</span><span class="sxs-lookup"><span data-stu-id="bfa8f-277">**Application Log:** No entry</span></span>

* <span data-ttu-id="bfa8f-278">**Journal stdout du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-278">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bfa8f-279">**Journal de débogage du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-279">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="bfa8f-280">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-280">Troubleshooting:</span></span>

<span data-ttu-id="bfa8f-281">Vérifiez que le pool d’applications n’est pas à l’état *Arrêté*.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-281">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="bfa8f-282">La sous-application inclut une section \<handlers></span><span class="sxs-lookup"><span data-stu-id="bfa8f-282">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="bfa8f-283">**Navigateur :** HTTP Erreur 500.19 - Erreur de serveur interne</span><span class="sxs-lookup"><span data-stu-id="bfa8f-283">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="bfa8f-284">**Journal des applications :** Entrée interdite</span><span class="sxs-lookup"><span data-stu-id="bfa8f-284">**Application Log:** No entry</span></span>

* <span data-ttu-id="bfa8f-285">**Journal stdout du module ASP.NET Core :** Le fichier journal de l’application racine est créé et indique un fonctionnement normal.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-285">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="bfa8f-286">Le fichier journal de la sous-application n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-286">The sub-app's log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bfa8f-287">**Journal de débogage du module ASP.NET Core :** Le fichier journal de l’application racine est créé et indique un fonctionnement normal.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-287">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="bfa8f-288">Le fichier journal de la sous-application n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-288">The sub-app's log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="bfa8f-289">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-289">Troubleshooting:</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="bfa8f-290">Vérifiez que le fichier *web.config* de la sous-application n’inclut pas de section `<handlers>` ou que la sous-application n’hérite pas des gestionnaires de l’application parente.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-290">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="bfa8f-291">La section `<system.webServer>` de l’application parente de *web.config* est placée à l’intérieur d’un élément `<location>`.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-291">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="bfa8f-292">La propriété <xref:System.Configuration.SectionInformation.InheritInChildApplications*> a la valeur `false` pour indiquer que les paramètres spécifiés dans l’élément [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) ne sont pas hérités par les applications situées dans un sous-répertoire de l’application parente.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-292">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="bfa8f-293">Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-293">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="bfa8f-294">Vérifiez que le fichier *web.config* de la sous-application n’inclut pas de section `<handlers>`.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-294">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

::: moniker-end

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="bfa8f-295">Chemin du journal stdout incorrect</span><span class="sxs-lookup"><span data-stu-id="bfa8f-295">stdout log path incorrect</span></span>

* <span data-ttu-id="bfa8f-296">**Navigateur :** L’application répond normalement.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-296">**Browser:** The app responds normally.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bfa8f-297">**Journal des applications :** Impossible de démarrer la redirection de stdout dans C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-297">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="bfa8f-298">Message d'exception : HRESULT 0x80070005 retourné sur {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-298">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="bfa8f-299">Impossible d’arrêter la redirection de stdout dans C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-299">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="bfa8f-300">Message d'exception : HRESULT 0x80070002 retourné sur {CHEMIN}.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-300">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="bfa8f-301">Impossible de démarrer la redirection de stdout dans {CHEMIN}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-301">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="bfa8f-302">**Journal stdout du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-302">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="bfa8f-303">**Journal de débogage du module ASP.NET Core :** Impossible de démarrer la redirection de stdout dans C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-303">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="bfa8f-304">Message d'exception : HRESULT 0x80070005 retourné sur {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-304">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="bfa8f-305">Impossible d’arrêter la redirection de stdout dans C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-305">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="bfa8f-306">Message d'exception : HRESULT 0x80070002 retourné sur {CHEMIN}.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-306">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="bfa8f-307">Impossible de démarrer la redirection de stdout dans {CHEMIN}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-307">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="bfa8f-308">**Journal des applications :** Avertissement : Impossible de créer stdoutLogFile \\?\{CHEMIN}\path_doesnt_exist\stdout_{ID_DE_PROCESSUS}_{HORODATAGE}.log. Code d’erreur = -2147024893.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-308">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="bfa8f-309">**Journal stdout du module ASP.NET Core :** Le fichier journal n’est pas créé.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-309">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="bfa8f-310">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-310">Troubleshooting:</span></span>

* <span data-ttu-id="bfa8f-311">Le chemin `stdoutLogFile` spécifié dans l’élément `<aspNetCore>` de *web.config* n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-311">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="bfa8f-312">Pour plus d’informations, consultez [Module ASP.NET Core : Création et redirection de journal](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span><span class="sxs-lookup"><span data-stu-id="bfa8f-312">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="bfa8f-313">L’utilisateur du pool d’applications ne dispose pas d’un accès en écriture sur le chemin du journal stdout.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-313">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="bfa8f-314">Problème général lié à la configuration d’application</span><span class="sxs-lookup"><span data-stu-id="bfa8f-314">Application configuration general issue</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="bfa8f-315">**Navigateur :** Erreur HTTP 500.0 - Échec du chargement du gestionnaire in-process d’ANCM **--OU--** Erreur HTTP 500.30 - Échec du démarrage in-process d’ANCM</span><span class="sxs-lookup"><span data-stu-id="bfa8f-315">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="bfa8f-316">**Journal des applications :** Variable</span><span class="sxs-lookup"><span data-stu-id="bfa8f-316">**Application Log:** Variable</span></span>

* <span data-ttu-id="bfa8f-317">**Journal stdout du module ASP.NET Core :** Le fichier journal est créé mais il est vide ou a été créé avec des entrées normales jusqu’au point d’échec de l’application.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-317">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="bfa8f-318">**Journal de débogage du module ASP.NET Core :** Variable</span><span class="sxs-lookup"><span data-stu-id="bfa8f-318">**ASP.NET Core Module Debug Log:** Variable</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="bfa8f-319">**Navigateur :** Erreur HTTP 502.5 - Échec du processus</span><span class="sxs-lookup"><span data-stu-id="bfa8f-319">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="bfa8f-320">**Journal des applications :** L’application « MACHINE/WEBROOT/APPHOST/{ASSEMBLY} » ayant pour racine physique « C:\{CHEMIN}\' » a créé un processus à l’aide de la ligne de commande « C:\{CHEMIN}\{ASSEMBLY}.{exe|dll} ». Toutefois, l’application a planté, n’a pas répondu ou n’a pas écouté sur le port indiqué « {PORT} ». Code d’erreur = « {CODE D’ERREUR} »</span><span class="sxs-lookup"><span data-stu-id="bfa8f-320">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="bfa8f-321">**Journal stdout du module ASP.NET Core :** Le fichier journal est créé mais il est vide.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-321">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="bfa8f-322">Résolution des problèmes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-322">Troubleshooting:</span></span>

<span data-ttu-id="bfa8f-323">Le processus n’a pas pu démarrer, probablement en raison d’un problème de configuration ou de programmation d’application.</span><span class="sxs-lookup"><span data-stu-id="bfa8f-323">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="bfa8f-324">Pour plus d’informations, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="bfa8f-324">For more information, see the following topics:</span></span>

* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:test/troubleshoot>
