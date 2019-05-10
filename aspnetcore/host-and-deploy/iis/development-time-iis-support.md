---
title: Prise en charge d’IIS pendant le développement dans Visual Studio pour ASP.NET Core
author: guardrex
description: Découvrez la prise en charge du débogage des applications ASP.NET Core en cas d’exécution avec IIS sur Windows Server.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: d2b2456c7ab6b72f2270b6edc17000695061cc2b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887754"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="9f6bc-103">Prise en charge d’IIS pendant le développement dans Visual Studio pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f6bc-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="9f6bc-104">De [Sourabh Shirhatti](https://twitter.com/sshirhatti) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9f6bc-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9f6bc-105">Cet article décrit la prise en charge de [Visual Studio](https://visualstudio.microsoft.com) pour le débogage des applications ASP.NET Core s’exécutant avec IIS sur Windows Server.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-105">This article describes [Visual Studio](https://visualstudio.microsoft.com) support for debugging ASP.NET Core apps running with IIS on Windows Server.</span></span> <span data-ttu-id="9f6bc-106">Cette rubrique présente ce scénario et la configuration d’un projet.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-106">This topic walks through enabling this scenario and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9f6bc-107">Prérequis</span><span class="sxs-lookup"><span data-stu-id="9f6bc-107">Prerequisites</span></span>

* [<span data-ttu-id="9f6bc-108">Visual Studio pour Windows</span><span class="sxs-lookup"><span data-stu-id="9f6bc-108">Visual Studio for Windows</span></span>](https://visualstudio.microsoft.com/downloads/)
* <span data-ttu-id="9f6bc-109">Charge de travail **Développement web et ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="9f6bc-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="9f6bc-110">Charge de travail **Développement multiplateforme .NET Core**</span><span class="sxs-lookup"><span data-stu-id="9f6bc-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="9f6bc-111">Certificat de sécurité X.509 (pour la prise en charge HTTPS)</span><span class="sxs-lookup"><span data-stu-id="9f6bc-111">X.509 security certificate (for HTTPS support)</span></span>

## <a name="enable-iis"></a><span data-ttu-id="9f6bc-112">Activer IIS</span><span class="sxs-lookup"><span data-stu-id="9f6bc-112">Enable IIS</span></span>

1. <span data-ttu-id="9f6bc-113">Dans Windows, accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran).</span><span class="sxs-lookup"><span data-stu-id="9f6bc-113">In Windows, navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="9f6bc-114">Cochez la case **Services IIS (Internet Information Services)**.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-114">Select the **Internet Information Services** check box.</span></span> <span data-ttu-id="9f6bc-115">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-115">Select **OK**.</span></span>

<span data-ttu-id="9f6bc-116">L’installation d’IIS peut nécessiter un redémarrage du système.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="9f6bc-117">Configurer IIS</span><span class="sxs-lookup"><span data-stu-id="9f6bc-117">Configure IIS</span></span>

<span data-ttu-id="9f6bc-118">IIS doit avoir un site web configuré avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="9f6bc-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="9f6bc-119">**Nom d’hôte** &ndash; C’est en règle générale le **Site web par défaut** qui est utilisé avec le **Nom d’hôte** `localhost`.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-119">**Host name** &ndash; Typically, the **Default Web Site** is used with a **Host name** of `localhost`.</span></span> <span data-ttu-id="9f6bc-120">Toutefois, n’importe quel site web IIS valide avec un nom d’hôte unique fonctionnera.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-120">However, any valid IIS website with a unique host name works.</span></span>
* <span data-ttu-id="9f6bc-121">**Liaison de site**</span><span class="sxs-lookup"><span data-stu-id="9f6bc-121">**Site Binding**</span></span>
  * <span data-ttu-id="9f6bc-122">Pour les applications qui exigent le protocole HTTPS, créez une liaison au port 443 avec un certificat.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-122">For apps that require HTTPS, create a binding to port 443 with a certificate.</span></span> <span data-ttu-id="9f6bc-123">On utilise en général le **certificat de développement IIS Express**, mais tous les certificats valides conviennent.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-123">Typically, the **IIS Express Development Certificate** is used, but any valid certificate works.</span></span>
  * <span data-ttu-id="9f6bc-124">Pour les applications qui utilisent le protocole HTTP, vérifiez l’existence d’une liaison au port 80 ou créez-en une pour un nouveau site.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-124">For apps that use HTTP, confirm the existence of a binding to post 80 or create a binding to port 80 for a new site.</span></span>
  * <span data-ttu-id="9f6bc-125">Utilisez une liaison unique pour HTTP ou HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-125">Use a single binding for either HTTP or HTTPS.</span></span> <span data-ttu-id="9f6bc-126">**La liaison simultanée aux ports HTTP et aux ports HTTPS n’est pas prise en charge.**</span><span class="sxs-lookup"><span data-stu-id="9f6bc-126">**Binding to both HTTP and HTTPS ports simultaneously isn't supported.**</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="9f6bc-127">Activer la prise en charge d’IIS pendant le développement dans Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f6bc-127">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="9f6bc-128">Lancez le programme d’installation de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-128">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="9f6bc-129">Sélectionnez **Modifier** pour l’installation de Visual Studio que vous souhaitez utiliser dans le cadre de la prise en charge IIS lors du développement.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-129">Select **Modify** for the Visual Studio installation that you plan to use for IIS development-time support.</span></span>
1. <span data-ttu-id="9f6bc-130">Pour la charge de travail **ASP.NET et développement web**, recherchez et installez le composant **Prise en charge IIS lors du développement**.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-130">For the **ASP.NET and web development** workload, locate and install the **Development time IIS support** component.</span></span>

   <span data-ttu-id="9f6bc-131">Le composant se trouve dans la section **Facultatif**, sous **Prise en charge IIS lors du développement**, dans le volet **Détails de l’installation** à droite des charges de travail.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-131">The component is listed in the **Optional** section under **Development time IIS support** in the **Installation details** panel to the right of the workloads.</span></span> <span data-ttu-id="9f6bc-132">Ce composant installe le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module), qui est un module IIS natif nécessaire à l’exécution des applications ASP.NET Core avec IIS.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-132">The component installs the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="9f6bc-133">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="9f6bc-133">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="9f6bc-134">Redirection HTTPS</span><span class="sxs-lookup"><span data-stu-id="9f6bc-134">HTTPS redirection</span></span>

<span data-ttu-id="9f6bc-135">Pour un nouveau projet qui exige le protocole HTTPS, cochez la case **Configurer pour HTTPS** dans la fenêtre **Créer une application web ASP.NET Core**</span><span class="sxs-lookup"><span data-stu-id="9f6bc-135">For a new project that requires HTTPS, select the check box to **Configure for HTTPS** in the **Create a new ASP.NET Core Web Application** window.</span></span> <span data-ttu-id="9f6bc-136">afin d’ajouter [Redirection HTTPS et middleware HSTS](xref:security/enforcing-ssl) à l’application lors de sa création.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-136">Selecting the check box adds [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) to the app when it's created.</span></span>

<span data-ttu-id="9f6bc-137">Pour un projet existant qui exige le protocole HTTPS, utilisez Redirection HTTPS et middleware HSTS dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-137">For an existing project that requires HTTPS, use HTTPS Redirection and HSTS Middleware in `Startup.Configure`.</span></span> <span data-ttu-id="9f6bc-138">Pour plus d'informations, consultez <xref:security/enforcing-ssl>.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-138">For more information, see <xref:security/enforcing-ssl>.</span></span>

<span data-ttu-id="9f6bc-139">Pour un projet qui utilise le protocole HTTP, [Redirection HTTPS et middleware HSTS](xref:security/enforcing-ssl) ne sont pas ajoutés à l’application.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-139">For a project that uses HTTP, [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) aren't added to the app.</span></span> <span data-ttu-id="9f6bc-140">Aucune configuration de l’application n’est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-140">No app configuration is required.</span></span>

### <a name="iis-launch-profile"></a><span data-ttu-id="9f6bc-141">Profil de lancement IIS</span><span class="sxs-lookup"><span data-stu-id="9f6bc-141">IIS launch profile</span></span>

<span data-ttu-id="9f6bc-142">Créez un profil de lancement pour ajouter la prise en charge d’IIS pendant le développement :</span><span class="sxs-lookup"><span data-stu-id="9f6bc-142">Create a new launch profile to add development-time IIS support:</span></span>

::: moniker range=">= aspnetcore-3.0"

1. <span data-ttu-id="9f6bc-143">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-143">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="9f6bc-144">Sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-144">Select **Properties**.</span></span> <span data-ttu-id="9f6bc-145">Ouvrez l’onglet **Déboguer**.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-145">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="9f6bc-146">Pour **Profil**, sélectionnez le bouton **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-146">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="9f6bc-147">Nommez le profil « IIS » dans la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-147">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="9f6bc-148">Sélectionnez **OK** pour créer le profil.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-148">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="9f6bc-149">Pour le paramètre **Lancer**, sélectionnez **IIS** dans la liste.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-149">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="9f6bc-150">Cochez la case **Lancer le navigateur** et indiquez l’URL du point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-150">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="9f6bc-151">Lorsque le protocole HTTPS est requis par l’application, utilisez un point de terminaison HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="9f6bc-151">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="9f6bc-152">Pour HTTP, utilisez un point de terminaison HTTP (`http://`).</span><span class="sxs-lookup"><span data-stu-id="9f6bc-152">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="9f6bc-153">Indiquez le nom d’hôte et le port utilisés par la [configuration IIS spécifiée précédemment](#configure-iis), en général `localhost`.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-153">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="9f6bc-154">Indiquez le nom de l’application à la fin de l’URL.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-154">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="9f6bc-155">Par exemple, `https://localhost/WebApplication1` (HTTPS) ou `http://localhost/WebApplication1` (HTTP) sont des URL de point de terminaison valide.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-155">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="9f6bc-156">Dans la section **Variables d’environnement**, sélectionnez le bouton **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-156">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="9f6bc-157">Indiquez une variable d’environnement ayant pour **Nom** `ASPNETCORE_ENVIRONMENT` et pour **Valeur** `Development`.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-157">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="9f6bc-158">Dans la zone **Paramètres de serveur web**, donnez à **URL de l’application** la valeur utilisée pour l’URL du point de terminaison **Lancer le navigateur**.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-158">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="9f6bc-159">Pour le paramètre **Modèle d’hébergement** dans Visual Studio 2019 (ou version ultérieure), sélectionnez **Par défaut** afin d’utiliser le modèle d’hébergement du projet.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-159">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="9f6bc-160">C’est la valeur de la propriété `<AspNetCoreHostingModel>` (`InProcess` ou `OutOfProcess`) qui est employée si elle est définie par le projet dans son fichier de projet.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-160">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="9f6bc-161">En l’absence de cette propriété, le modèle d’hébergement par défaut de l’application est utilisé in-process.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-161">If the property isn't present, the default hosting model of the app is used, which is in-process.</span></span> <span data-ttu-id="9f6bc-162">Si l’application réclame un paramètre de modèle d’hébergement explicite autre que son modèle normal, définissez le **Modèle d’hébergement** sur `In Process` ou `Out Of Process` en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-162">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="9f6bc-163">Enregistrez le profil.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-163">Save the profile.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

1. <span data-ttu-id="9f6bc-164">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-164">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="9f6bc-165">Sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-165">Select **Properties**.</span></span> <span data-ttu-id="9f6bc-166">Ouvrez l’onglet **Déboguer**.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-166">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="9f6bc-167">Pour **Profil**, sélectionnez le bouton **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-167">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="9f6bc-168">Nommez le profil « IIS » dans la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-168">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="9f6bc-169">Sélectionnez **OK** pour créer le profil.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-169">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="9f6bc-170">Pour le paramètre **Lancer**, sélectionnez **IIS** dans la liste.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-170">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="9f6bc-171">Cochez la case **Lancer le navigateur** et indiquez l’URL du point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-171">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="9f6bc-172">Lorsque le protocole HTTPS est requis par l’application, utilisez un point de terminaison HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="9f6bc-172">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="9f6bc-173">Pour HTTP, utilisez un point de terminaison HTTP (`http://`).</span><span class="sxs-lookup"><span data-stu-id="9f6bc-173">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="9f6bc-174">Indiquez le nom d’hôte et le port utilisés par la [configuration IIS spécifiée précédemment](#configure-iis), en général `localhost`.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-174">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="9f6bc-175">Indiquez le nom de l’application à la fin de l’URL.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-175">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="9f6bc-176">Par exemple, `https://localhost/WebApplication1` (HTTPS) ou `http://localhost/WebApplication1` (HTTP) sont des URL de point de terminaison valide.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-176">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="9f6bc-177">Dans la section **Variables d’environnement**, sélectionnez le bouton **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-177">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="9f6bc-178">Indiquez une variable d’environnement ayant pour **Nom** `ASPNETCORE_ENVIRONMENT` et pour **Valeur** `Development`.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-178">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="9f6bc-179">Dans la zone **Paramètres de serveur web**, donnez à **URL de l’application** la valeur utilisée pour l’URL du point de terminaison **Lancer le navigateur**.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-179">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="9f6bc-180">Pour le paramètre **Modèle d’hébergement** dans Visual Studio 2019 (ou version ultérieure), sélectionnez **Par défaut** afin d’utiliser le modèle d’hébergement du projet.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-180">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="9f6bc-181">C’est la valeur de la propriété `<AspNetCoreHostingModel>` (`InProcess` ou `OutOfProcess`) qui est employée si elle est définie par le projet dans son fichier de projet.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-181">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="9f6bc-182">En l’absence de cette propriété, le modèle d’hébergement par défaut de l’application est utilisé hors processus.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-182">If the property isn't present, the default hosting model of the app is used, which is out-of-process.</span></span> <span data-ttu-id="9f6bc-183">Si l’application réclame un paramètre de modèle d’hébergement explicite autre que son modèle normal, définissez le **Modèle d’hébergement** sur `In Process` ou `Out Of Process` en fonction des besoins.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-183">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="9f6bc-184">Enregistrez le profil.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-184">Save the profile.</span></span>

::: moniker-end

<span data-ttu-id="9f6bc-185">Si vous n’utilisez pas Visual Studio, ajoutez manuellement un profil de lancement au fichier [launchSettings.json](http://json.schemastore.org/launchsettings) dans le dossier *Propriétés*.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-185">When not using Visual Studio, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the *Properties* folder.</span></span> <span data-ttu-id="9f6bc-186">L’exemple suivant configure le profil de façon à utiliser le protocole HTTPS :</span><span class="sxs-lookup"><span data-stu-id="9f6bc-186">The following example configures the profile to use the HTTPS protocol:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

<span data-ttu-id="9f6bc-187">Vérifiez que les points de terminaison `applicationUrl` et `launchUrl` coïncident et utilisent le même protocole que la configuration de liaison IIS, c’est-à-dire HTTP ou HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-187">Confirm that the `applicationUrl` and `launchUrl` endpoints match and use the same protocol as the IIS binding configuration, either HTTP or HTTPS.</span></span>

## <a name="run-the-project"></a><span data-ttu-id="9f6bc-188">Exécuter le projet</span><span class="sxs-lookup"><span data-stu-id="9f6bc-188">Run the project</span></span>

<span data-ttu-id="9f6bc-189">Exécutez Visual Studio en tant qu’administrateur :</span><span class="sxs-lookup"><span data-stu-id="9f6bc-189">Run Visual Studio as an administrator:</span></span>

* <span data-ttu-id="9f6bc-190">Vérifiez que la liste déroulante des configurations de build est définie sur **Déboguer**.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-190">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="9f6bc-191">Définissez le bouton Exécuter sur le profil **IIS** et sélectionnez le bouton pour démarrer l’application.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-191">Set the Run button to the **IIS** profile and select the button to start the app.</span></span>

<span data-ttu-id="9f6bc-192">Visual Studio peut demander un redémarrage si vous ne l’exécutez pas en tant qu’administrateur.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-192">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="9f6bc-193">Si vous y êtes invité, redémarrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-193">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="9f6bc-194">Si vous utilisez un certificat de développement non approuvé, le navigateur peut vous amener à créer une exception pour ce certificat.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-194">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="9f6bc-195">Le débogage d’une configuration de build de mise en production avec [Uniquement mon code](/visualstudio/debugger/just-my-code) et des optimisations du compilateur entraîne une baisse des résultats.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-195">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="9f6bc-196">Par exemple, les points d’arrêt ne sont pas atteints.</span><span class="sxs-lookup"><span data-stu-id="9f6bc-196">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9f6bc-197">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9f6bc-197">Additional resources</span></span>

* [<span data-ttu-id="9f6bc-198">Bien démarrer avec le Gestionnaire IIS dans IIS</span><span class="sxs-lookup"><span data-stu-id="9f6bc-198">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="9f6bc-199">Héberger ASP.NET Core sur Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="9f6bc-199">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="9f6bc-200">Introduction au Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f6bc-200">Introduction to ASP.NET Core Module</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="9f6bc-201">Informations de référence sur la configuration du Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f6bc-201">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="9f6bc-202">Appliquer HTTPS</span><span class="sxs-lookup"><span data-stu-id="9f6bc-202">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
